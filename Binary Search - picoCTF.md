Here is the writeup for "Binary Search" a Easy-level CTF challenge that is categorized as a General Skill, without further ado let's get into it!

First it will give us the intro to the challenge and what to do to get us started.
![image](https://github.com/user-attachments/assets/5d5d926a-e002-4e09-9006-0b8b442d6d14)
The challenge here refers to the concept of binary search as an important algorithm used to quickly find an item in a sorted list. Here, you need to locate 
a "flag" hidden in 1,000 possible cases but with only 10 guesses available to reach a successful conclusion-you know it's a case for binary search.

Cybersecurity experts frequently cope with volumes of data-logs, vulnerability reports, and forensic artifacts. This challenge would develop in you 
the ability to conduct a binary search but also stress the importance of employing highly efficient methods while dealing with data.

So let's start doing the challenge, we can first download the file that was given, which was challenge.zip, and the contents is a .sh file that makes it
a Bash script that facilitates the guessing process or provides the interface for applying the binary search algorithm. After that I opened the script, which revealed
this code:


            #!/bin/bash

            # Generate a random number between 1 and 1000
            target=$(( (RANDOM % 1000) + 1 ))

            echo "Welcome to the Binary Search Game!"
            echo "I'm thinking of a number between 1 and 1000."

            # Trap signals to prevent exiting
            trap 'echo "Exiting is not allowed."' INT
            trap '' SIGQUIT
            trap '' SIGTSTP

            # Limit the player to 10 guesses
            MAX_GUESSES=10
            guess_count=0

            while (( guess_count < MAX_GUESSES )); do
                read -p "Enter your guess: " guess

                if ! [[ "$guess" =~ ^[0-9]+$ ]]; then
                    echo "Please enter a valid number."
                    continue
                fi

                (( guess_count++ ))

                if (( guess < target )); then
                    echo "Higher! Try again."
                elif (( guess > target )); then
                    echo "Lower! Try again."
                else
                    echo "Congratulations! You guessed the correct number: $target"

                    # Retrieve the flag from the metadata file
                    flag=$(cat /challenge/metadata.json | jq -r '.flag')
                    echo "Here's your flag: $flag"
                    exit 0  # Exit with success code
                fi
            done

            # Player has exceeded maximum guesses
            echo "Sorry, you've exceeded the maximum number of guesses."
            exit 1  # Exit with error code to close the connection

This code is basically doing the things that we we're told in the introduction, which is we need to guess a number between 1-1000, so let's do it.
We first need to ssh into the server using: ssh -p 65140 ctf-player@atlas.picoctf.net
Then we're greeted by this
![image](https://github.com/user-attachments/assets/7632f67e-d566-4d43-ae7b-21fc3be209cf)

We need to guess a number between 1 and 1000, and as the challenge title suggests, we’ll use binary search to solve it. This involves repeatedly splitting 
the range in half to narrow down the possibilities. Starting with 1000 options, we first guess 500. Based on the feedback ("higher" or "lower"), we then halve 
the remaining range, guessing either 250 or 750, and continue this process until we find the correct number.
![image](https://github.com/user-attachments/assets/629c47d2-d29b-4c39-97ad-4016e8b0031a)

As we can see by doing Binary Search I've successfully obtained the flag, which is picoCTF{g00d_gu355_ee8225d0}

That's all thank you for reading!
