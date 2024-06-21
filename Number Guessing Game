#!/bin/bash
PSQL="psql --username=freecodecamp --dbname=number_guess -t --no-align -c"

#prompt enter username
echo "Enter your username:"
read USERNAME

#username available
USERNAME_ID=$($PSQL "SELECT user_id FROM users WHERE username = '$USERNAME'")
#games played
GAMES_PLAYED=$($PSQL "SELECT COUNT(*) FROM users INNER JOIN games USING(user_id) WHERE username = '$USERNAME'")
#best game
BEST_GAME=$($PSQL "SELECT MIN(number_guesses) FROM users INNER JOIN games USING(user_id) WHERE username = '$USERNAME'")

#if username already exists
if [[ -z $USERNAME_ID ]]
  then
    INSERT_USERNAME=$($PSQL "INSERT INTO users(username) VALUES('$USERNAME')")
    echo "Welcome, $USERNAME! It looks like this is your first time here."
  else
    echo "Welcome back, $USERNAME! You have played $GAMES_PLAYED games, and your best game took $BEST_GAME guesses."
fi

#select random number
RANDOM_NUMBER=$((1+RANDOM % 1000))

#set guess variable
GUESS=1

#prompt for number guess
echo "Guess the secret number between 1 and 1000:"
while read NUM
  do
    if [[ ! $NUM =~ ^[0-9]+$ ]]
      then
        echo "That is not an integer, guess again:"
      else
        #guess is correct
        if [[ $NUM -eq $RANDOM_NUMBER ]]
          then
            break;
          else
            #if guess is too high
            if [[ $NUM -gt $RANDOM_NUMBER ]]
              then
                echo "It's lower than that, guess again:"
              elif [[ $NUM -lt $RANDOM_NUMBER ]]
                then
                  echo "It's higher than that, guess again:"
            fi 
        fi    
    fi
    GUESS=$(( $GUESS + 1 ))
done

  if [[ $GUESS == 1 ]]
  then
    echo "You guessed it in $GUESS tries. The secret number was $RANDOM_NUMBER. Nice job!"
  else
    echo "You guessed it in $GUESS tries. The secret number was $RANDOM_NUMBER. Nice job!"
  fi


#insert values
USER_ID=$($PSQL "SELECT user_id FROM users WHERE username = '$USERNAME'")
UPDATE_GAME=$($PSQL "INSERT INTO games(number_guesses, user_id) VALUES($GUESS, $USER_ID)")


