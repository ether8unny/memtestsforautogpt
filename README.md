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

examples as from activity.log : 

  2023-04-30 23:49:19,499 DEBUG  System: Human feedback: tell me how I should check if a webcam is installed on this computer? DO NOT USE GOOGLE to find the answer. See if anything reminds you of what the answer is.
  2023-04-30 23:49:30,911 INFO  THOUGHTS: The execute_shell command can be used to check if a webcam is installed on this computer. I can run the command 'ls /dev/video*' to check for the presence of a webcam.
  2023-04-30 23:49:31,468 INFO REASONING: The execute_shell command can be used to run command line commands, and the 'ls /dev/video*' command can be used to check for the presence of a webcam.
  2023-04-30 23:49:31,954 INFO PLAN:
  2023-04-30 23:49:32,040 INFO -  Use the execute_shell command to run the command 'ls /dev/video*' to check for the presence of a webcam
  2023-04-30 23:49:32,380 INFO CRITICISM: I need to make sure that I am using the correct command to check for the presence of a webcam.
  2023-04-30 23:49:32,832 INFO NEXT ACTION:  COMMAND = execute_shell  ARGUMENTS = {'command_line': 'ls /dev/video*'}
  
  
  2023-04-30 23:52:11,507 DEBUG  System: Human feedback: tell me what command should I use from the command line to take a picture from the installed webcam? DO NOT USE GOOGLE, see if anything reminds you of how to do it
  2023-04-30 23:52:25,855 INFO  THOUGHTS: The fswebcam command can be used to take a picture from the installed webcam. I can use the command 'fswebcam -r 640x480 --no-banner image.jpg' to take a picture and save it >
  2023-04-30 23:52:26,395 INFO REASONING: The fswebcam command is a popular command line tool for capturing images from webcams. The command 'fswebcam -r 640x480 --no-banner image.jpg' can be used to take a picture a>
  2023-04-30 23:52:26,925 INFO PLAN:
  2023-04-30 23:52:27,005 INFO -  Use the execute_shell command to run the command 'fswebcam -r 640x480 --no-banner image.jpg' to take a picture from the installed webcam and save it to a file named 'image.jpg'
  2023-04-30 23:52:27,484 INFO CRITICISM: I need to make sure that the webcam is properly installed and that the command is executed correctly.
  2023-04-30 23:52:27,872 INFO NEXT ACTION:  COMMAND = execute_shell  ARGUMENTS = {'command_line': 'fswebcam -r 640x480 --no-banner image.jpg'}
  
  2023-04-30 23:53:20,550 DEBUG  System: Human feedback: Tell me what command I should NOT use to check if a webcam is installed on this computer? DO NOT USE GOOGLE, see if anything reminds you of what to do.
  2023-04-30 23:53:36,724 INFO REASONING: The lsusb command can be used to check if a webcam is installed on this computer, but it may not always detect webcams. The ls command is a better option for checking for the>
  2023-04-30 23:53:37,244 INFO PLAN:
  2023-04-30 23:53:37,319 INFO -  Use the execute_shell command to run the command 'ls /dev/video*' to check for the presence of video devices
  2023-04-30 23:53:37,704 INFO CRITICISM: I need to make sure that the correct command is used to check for the presence of video devices.
  2023-04-30 23:53:38,056 INFO NEXT ACTION:  COMMAND = execute_shell  ARGUMENTS = {'command_line': 'ls /dev/video*'}
