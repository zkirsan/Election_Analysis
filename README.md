# Election_Analysis
## Project Overview
To finish the election audit of a recent local congressional election, a Colorado Board of Elections employee asked me the following jobs;

  1. Calculate the total number of votes cast.
  2. Get a complete list of candidates who received votes.
  3. Calculate the total number of votes each candidate received.
  4. Calculate the percentage of votes each candidate won.
  5. Determine the winner of the election based on the popular vote.

## Resources
  - Data Source: election_results.csv
  - Software: Python 3.7.6, Visual Studio Code, 1.54
## Summary
The analysis of the election shows that;
  - There were 369,711 votes cast in the election.
  - The candidates were;
      - Charles Casper Stockham
      - Diana DeGette
      - Raymon Anthony Doane
  - The candidate results were;
      - Charles Casper Stockham received 23.00% of the vote and 85,213 number of votes.
      - Diana DeGette received 73.8% of the vote and 272,892 number of votes.
      - Raymon Anthony Doane received 3.1% of the vote and 11,606 number of votes.
  - The winner of the election was;
      - Diana DeGette, who received 73.8% of the vote and 272,892 number of votes.

## Challenge Overview
The challenge was consisting of two technical analysis deliverables, which were;
  1. The Election Results Printed to the Command Line
  2. The Election Results Saved to a Text File
<p align="center"><img src="https://github.com/zkirsan/Election_Analysis/blob/main/Terminal_Record.PNG"></img></p>

## Challenge Summary

# -*- coding: UTF-8 -*-
"""PyPoll Homework Challenge Solution."""

# Add our dependencies.
import csv
import os

# Add a variable to load a file from a path.
file_to_load = os.path.join("Resources", "election_results.csv")
# Add a variable to save the file to a path.
file_to_save = os.path.join("analysis", "election_analysis.txt")


# Initialize a total vote counter.
total_votes = 0

# Candidate Options and candidate votes.
candidate_options = []
candidate_votes = {}

# 1: Create a county list and county votes dictionary.
county_options= []
county_votes={}


# Track the winning candidate, vote count and percentage
winning_candidate = ""
winning_count = 0
winning_percentage = 0

# 2: Track the largest county and county voter turnout.
largest_county = ""
largest_county_count= 0
largest_county_percentage=0

# Read the csv and convert it into a list of dictionaries
with open(file_to_load) as election_data:
    reader = csv.reader(election_data)

    # Read the header
    header = next(reader)

    # For each row in the CSV file.
    for row in reader:

        # Add to the total vote count
        total_votes = total_votes + 1

        # Get the candidate name from each row.
        candidate_name = row[2]

        # 3: Extract the county name from each row.
        county_name=row[1]

        # If the candidate does not match any existing candidate add it to
        # the candidate list
        if candidate_name not in candidate_options:

            # Add the candidate name to the candidate list.
            candidate_options.append(candidate_name)

            # And begin tracking that candidate's voter count.
            candidate_votes[candidate_name] = 0

        # Add a vote to that candidate's count
        candidate_votes[candidate_name] += 1

        # 4a: Write an if statement that checks that the
        # county does not match any existing county in the county list.
        if county_name not in county_options:

            # 4b: Add the existing county to the list of counties.
            county_options.append(county_name)

            # 4c: Begin tracking the county's vote count.
            county_votes[county_name]=0


        # 5: Add a vote to that county's vote count.
        county_votes[county_name] += 1
        


# Save the results to our text file.

with open(file_to_save, "w") as txt_file:

    # Print the final vote count (to terminal)
    election_results = (
        f"\nElection Results\n"
        f"-------------------------\n"
        f"Total Votes: {total_votes:,}\n"
        f"-------------------------\n"
        f"County Votes:\n")
    print(election_results, end="")

    txt_file.write(election_results)

    # 6a: Write a for loop to get the county from the county dictionary.
    for county in county_votes:

        # 6b: Retrieve the county vote count.
        votes=county_votes[county]

        # 6c: Calculate the percentage of votes for the county.
        vote_percentage= float(votes)/float(total_votes)*100


         # 6d: Print the county results to the terminal.
        county_results = ( f"{county}: {vote_percentage:.1f}% ({votes:,})\n")
        print(county_results)

         # 6e: Save the county votes to a text file.
        txt_file.write(county_results)
        

         # 6f: Write an if statement to determine the winning county and get its vote count.
        if (votes> largest_county_count) and (vote_percentage> largest_county_percentage):
            largest_county_count= votes
            largest_county_percentage = vote_percentage
            largest_county = county

    # 7: Print the county with the largest turnout to the terminal.
            largest_county_summary = (
                f"--------------------------\n"
                f"Largest County Turnout: {largest_county}\n"
                f"--------------------------\n")
    print(largest_county_summary)


    # 8: Save the county with the largest turnout to a text file.
    txt_file.write(largest_county_summary)

    # Save the final candidate vote count to the text file.
    for candidate_name in candidate_votes:

        # Retrieve vote count and percentage
        votes = candidate_votes.get(candidate_name)
        vote_percentage = float(votes) / float(total_votes) * 100
        candidate_results = (
            f"{candidate_name}: {vote_percentage:.1f}% ({votes:,})\n")

        # Print each candidate's voter count and percentage to the
        # terminal.
        print(candidate_results)
        #  Save the candidate results to our text file.
        txt_file.write(candidate_results)

        # Determine winning vote count, winning percentage, and candidate.
        if (votes > winning_count) and (vote_percentage > winning_percentage):
            winning_count = votes
            winning_percentage = vote_percentage
            winning_candidate = candidate_name

    # Print the winning candidate (to terminal)
    winning_candidate_summary = (
        f"-------------------------\n"
        f"Winner: {winning_candidate}\n"
        f"Winning Vote Count: {winning_count:,}\n"
        f"Winning Percentage: {winning_percentage:.1f}%\n"
        f"-------------------------\n")
    print(winning_candidate_summary)

    # Save the winning candidate's name to the text file
    txt_file.write(winning_candidate_summary)
