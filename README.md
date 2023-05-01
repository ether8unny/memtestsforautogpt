# memtestsforautogpt
results from doing contextual memory tests with autogpt


This is the result of memtesting autogpt 3.5turbo using local redis memory on stable-0.2.2

I'm including the .env, the full output from the console using debug, the associated logs, the init files I am using, i.e. memory and goals and important_info for the teaching run as well as the test run.

Summary of test :

On a prior instance I got autogpt to with fairly simple prompts taught it to check if a webcam is installed on my host machine, determine how to take a picture from it using CLI, save it, upload it to imgur and write the url to the goals.txt file. I used redis insight to examine the memories loaded into the database and extracted a set of memories that described both successes and failures in the process. I then trimmed them considerably into this format : 

"text": "I think I should start by determining if a webcam is installed on this computer."
Result: Command execute_shell "ls /dev/video*" returned: b'/dev/video0\n/dev/video1\n'b''. Success

skipping a line between memories. 

I then compiled them into a memory file with the intent of using the ingest function to add them to a freshly wiped database. The ingestion script was not functional so I instead opted to load the memories via a goals injection. Once the injection was complete I then quizzed the gent on how to correctly perfomr the tasks it previously used. I also asked which commands to NOT use for the tasks. The Agent was 100% correct in all cases on the first try. It was able to tell me both how to do a command successfully, as well as why i should not use other commands. 
