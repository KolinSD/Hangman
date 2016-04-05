# -*- coding: utf-8 -*-
# 6.00 Problem Set 3
# 
# Hangman game
#Irina Radosavljevic

# -----------------------------------
# Helper code
# You don't need to understand this helper code,
# but you will have to know how to use the functions
# (so be sure to read the docstrings!)

import random
import string

WORDLIST_FILENAME = "/Users/irina/Documents/6.00edX files/words.txt"

def loadWords():
    """
    Returns a list of valid words. Words are strings of lowercase letters.
    
    Depending on the size of the word list, this function may
    take a while to finish.
    """
    print "Loading word list from file..."
    # inFile: file
    inFile = open(WORDLIST_FILENAME, 'r', 0)
    # line: string
    line = inFile.readline()
    # wordlist: list of strings
    wordlist = string.split(line)
    print "  ", len(wordlist), "words loaded."
    return wordlist

def chooseWord(wordlist):
    """
    wordlist (list): list of words (strings)

    Returns a word from wordlist at random
    """
    return random.choice(wordlist)

# end of helper code
# -----------------------------------

# Load the list of words into the variable wordlist
# so that it can be accessed from anywhere in the program
wordlist = loadWords()


def isWordGuessed(secretWord, lettersGuessed):
    '''
    secretWord: string, the word the user is guessing
    lettersGuessed: list, what letters have been guessed so far
    returns: boolean, True if all the letters of secretWord are in lettersGuessed;
      False otherwise
    '''
    # FILL IN YOUR CODE HERE...
    s = str()
    for letter in secretWord:
        if letter in lettersGuessed:
            s += letter
    if s == secretWord:
        return True
    else:
        return False



def getGuessedWord(secretWord, lettersGuessed):
    '''
    secretWord: string, the word the user is guessing
    lettersGuessed: list, what letters have been guessed so far
    returns: string, comprised of letters and underscores that represents
      what letters in secretWord have been guessed so far.
    '''
    # FILL IN YOUR CODE HERE...
    # converting secretWord into a list with underscores instead of characters
    # each underscore belongs to certain index
    listB =[]
    for num in range (len(secretWord)):
        listB.append('_')
    
    
    # interation through the characters in secretWord
    for letter in secretWord:
        # test if this character has been guessed
        # is not, we check the next character
        if letter in lettersGuessed:
            # if so, then we check how much time these characters appear
            # at the secretWord
            if secretWord.count(letter) == 1:
                # if it is only one, we change our list with this letter
                # on the place of its index
                listB [secretWord.find(letter)] = letter
            # if it is more than one
            else:
                # we check the secretWord by steps
                # by slicing it
                i = 0
                while i < len(secretWord):
                    if secretWord.find(letter,i,i+1) != -1:
                        # if the character is in the slice, we change the list
                        # with this character on the place of its index
                        listB [secretWord.find(letter,i,i+1)] = letter
                    i += 1
                
     # converting list back to the string       
    return ''.join(listB)


def getAvailableLetters(lettersGuessed):
    '''
    lettersGuessed: list, what letters have been guessed so far
    returns: string, comprised of letters that represents what letters have not
      yet been guessed.
    '''
    # FILL IN YOUR CODE HERE...
    # importing module with strings functions to create a string with 
    #all alphabet
    import string
    # converting all alphabet string into the list to have ability to change it
    listC =[]
    for char in string.ascii_lowercase:
        listC.append(char)
     
    # removing guested letters from all alphabet list   
    for letter in lettersGuessed:
        if letter in listC:
            listC.remove(letter)
     
    # converting the result into a string       
    return ''.join(listC)
    

def hangman(secretWord):
    '''
    secretWord: string, the secret word to guess.

    Starts up an interactive game of Hangman.

    * At the start of the game, let the user know how many 
      letters the secretWord contains.

    * Ask the user to supply one guess (i.e. letter) per round.

    * The user should receive feedback immediately after each guess 
      about whether their guess appears in the computers word.

    * After each round, you should also display to the user the 
      partially guessed word so far, as well as letters that the 
      user has not yet guessed.

    Follows the other limitations detailed in the problem write-up.
    '''
    # FILL IN YOUR CODE HERE...
   
    print 'Welcome to the game, Hangman!'  # to start from the greeting
    # inform how many letter the word contains
    print  'I am thinking of a word that is ' + str(len(secretWord)) + ' letters long.'
    # the lin, divading parts for better usability
    print '_________'
            
            
    lettersGuessed = [] # the list containing all the letters user unputs
    mistakes = 0 # to count mistakes
    tries = 8 # to count tries
    
    
    while mistakes < 8:  # the rule of the game
        print 'You have '+ str(tries) + ' guesses left.' # inform the user how many tries 
        # he has left
        print 'Available letters: ' + getAvailableLetters(lettersGuessed)
        guess = raw_input('Please guess a letter: ')
        guessInLowerCase = guess.lower()# converting letter to lower
        guess = guessInLowerCase 

            
        if guess in lettersGuessed: # checks if user already used this letter and 
            # if he did, we ask to try again
            print "Oops! You've already guessed that letter: " + getGuessedWord(secretWord, lettersGuessed)
            print '_________'
            
        elif len(guess) > 1:
            print "Oops! You've entered more than one letter. Try again!" + getGuessedWord(secretWord, lettersGuessed)
            print '_________'
            
 
        # if guess is right               
        elif guess in secretWord:
            lettersGuessed += guess # we add letter to the letters guessed list
            print 'Good guess: ' + getGuessedWord(secretWord, lettersGuessed)
            print '_________'
 
            if isWordGuessed(secretWord, lettersGuessed) == True:# then we check if the world is guessed
                    print 'Congratulations, you won!'
                    print '_________'
                    break # if so we break the program
            
        
        else:
            print 'Oops! That letter is not in my word: ' + getGuessedWord(secretWord, lettersGuessed)
            mistakes += 1 # if it is a wrong guess, we count mistakes
            tries -= 1
            lettersGuessed += guess
            print '_________'

            
    else: # is user is out of tries, game is over
        print 'Sorry, you ran out of guesses. The word was else.'
        
# When you've completed your hangman function, uncomment these two lines
# and run this file to test! (hint: you might want to pick your own
# secretWord while you're testing)

secretWord = chooseWord(wordlist).lower()
hangman(secretWord)
