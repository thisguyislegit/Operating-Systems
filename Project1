//Kyle Bergeron
//CSCI 3341: Intro to Operating Systems
//Professor: Ray Hashemi
//Assignment 1
//DUE: October 11, 201

#include <iostream>
#include <pthread.h>
#include <fstream>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <vector>
#include <algorithm>
#include <ctype.h>
#include <map>

using namespace std;
void *sifterT (void *);
void *decoderT (void *);
void *fenceT (void *);
void *hillT (void *);
void *valleyT (void *);
bool isValidInput (string input);
string section1, section2, section3;

int
main ()
{
  string input;
  pthread_t sifter;

  //Sifter thread
  if (pthread_create (&sifter, NULL, sifterT, &input))
    {
      cout << "Error creating sifter thread";
      return 0;
    }

  if (pthread_join (sifter, NULL))
    {
      cout << "Error joining sifter thread";
      return 0;
    }

  return 0;
}

//Sifter Thread
void *
sifterT (void *in)
{
  string *input = (string *) (in);
  string message;
int i;
for (int i = 0; i < 3; i)
    {
      cout << "Enter your message to exit type (exit now): " << endl;
      getline (cin, message);
       //message = "***ABC 5 6 7 8 10 1 4 8 11 *K SAAPXGOW**MKPW 10 2 12 1*J RZZOWFNV";
       
      if (message == "exit now")
	{
	  cout << "You exited the program" << endl;
	  return EXIT_SUCCESS;
	}
      section1.clear ();
      section2.clear ();
      section3.clear ();


      //check if input is valid
      if (isValidInput (message))
	{

	  //if valid reset counter
	  i = 0;
	  cout << "Valid input was recieved starting decoder thread." << endl;

	  //start decoder thread
	  pthread_t decoder;

	  //check if decoder was successfully created
	  if (pthread_create (&decoder, nullptr, decoderT, &message))
	    {
	      cout << "Error creating the decode thread";
	      return 0;
	    }

	  //check if decoder was successfully joined
	  if (pthread_join (decoder, nullptr))
	    {
	      cout << "Error joining the decoder thread";
	      return 0;
	    }


	}
      else
	{
	  if (i == 0)
	    {
	      i += 1;
	      cout <<
		"Invalid Input you have two more tries to input a vaild message."
		<< endl;
	    }
	  else if (i == 1)
	    {
	      i += 1;
	      cout <<
		"Invalid Input you have one more try to input a valid message."
		<< endl;
	    }
	  else if (i == 2)
	    {
	      i += 1;
	      cout << "Invalid Input you have no more tries left." << endl;

	    }
	  else
	    {
	      cout << "There was an error.";
	    }
	  message.clear ();
	}
    }
  return EXIT_SUCCESS;

}

//Helper for Sifter: checks if each section can be found and identifies each section 
bool
isValidInput (string message)
{
  bool valid = true;
  bool section1Valid = false;
  bool section2Valid = false;
  bool section3Valid = false;

  // find where the * are
  int i = 0;
  while (i < message.length () - 1 && valid)
    {
      int asterCount = 0;

      //eliminate white space before asterics
      while (isspace(message[i])){
	i++;
      }

      //if there is one asteric count
      if (message[i] == '*')
	{
	  asterCount = 1;
	}

      //if the next two are asteric overide count
      if (message[i + 1] == '*')
	{
	  asterCount = 2;

	  //if the next three are asteric overide count
	}
      if (message[i + 2] == '*')
	{
	  asterCount = 3;
	}

//find asteric one message and check if asteric one is already done
      if (asterCount == 1)
	{
	  i++;
	  for (int j = i;
	       message[j] != '*' && i <= message.length () - 1
	       && !section1Valid; j++, i++)
	    {
	      section1 = section1 + message[j];
	    }

	  //if asteric one has already been done set to invalid message
	  if (section1Valid)
	    {
	      valid = false;
	    }

	  //set that asteric one is done
	  section1Valid = true;
	}

      //find asteric two message and chekc if asteric two is already done
      else if (asterCount == 2)
	{
	  i = i + 2;
	  for (int j = i;
	       message[j] != '*' && i <= message.length () - 1
	       && !section2Valid; j++, i++)
	    {
	      section2 = section2 + message[j];
	    }
	  //if asteric two has already been done set to invalid message
	  if (section2Valid)
	    {
	      valid = false;
	    }
	  //set that asteric two is done
	  section2Valid = true;
	}
      //find asteric three message and check if asteric three is already done
      else if (asterCount == 3 && !section3Valid)
	{
	  i = i + 3;
	  for (int j = i;
	       message[j] != '*' && i <= message.length () - 1; j++, i++)
	    {
	      section3 = section3 + message[j];
	    }
	  //if asteric three has already been done set to invalid message
	  if (section3Valid)
	    {
	      valid = false;
	    }
	  //set that asteric three is done
	  section3Valid = true;
	}
      else
	{
	  valid = false;
	}
    }

  //output sections before they are encoded/decoded
  cout << "Section 1: " << section1 << endl << "Section 2: " << section2
    << endl << "Section 3: " << section3 << endl;

  //check that the message is valid
  if (valid && section1Valid && section2Valid && section3Valid)
    {
      return true;
    }
  else
    {
      return false;
    }
}

//Decoder Thread
void *
decoderT (void *in)
{

  void *threadPointer;
  pthread_t fence, hill, valley;

     if (pthread_create(&fence, NULL, fenceT, &section1)) {
     cout << "Error creating Fence thread";
     return 0;
     }
     if (pthread_join(fence, NULL)) {
     cout << "Error joining Fence thread";
     return 0;
     }
     
  if (pthread_create (&hill, NULL, hillT, &section2))
    {
      cout << "Error creating Hill thread";
      return 0;
    }

  if (pthread_join (hill, &threadPointer))
    {
      cout << "Error joining Hill thread" << threadPointer;
      return 0;
    }


     if (pthread_create(&valley, NULL, valleyT, &section3)) 
     {
     cout << "Error creating Valley thread";
     return 0;
     }

     if (pthread_join(valley, NULL)) {
     cout << "Error joining Valley thread";
     return 0;
     }
   
  return EXIT_SUCCESS;
}

//Algorithm #1: Rail Fence Algorithm
void *
fenceT (void *in)
{
  string *input = (string *) (in);
  string message = *input;
  string encryptedMessage;
  string section1FPart1;
  string section1FPart2;
  string section2F;
  int n =-1;
  int j;
  int max;
  bool section2FFound=true;

//read whitespace before section1F
  int i = 0;
  while (isspace (message[i]))
    {
      i++;
    }

  //get the message for section1F and section2F
 for (i; i < message.length () && section2FFound; i++){
      
    if (isspace(message[i])){
	    //read space
	}
    else if ( isdigit (message[i]))
	{
	 section1FPart1+=message[i];
	}
    else if ( isalpha(message[i]))
	{
	  	  section2F = message.substr(i, string::npos);
	  	  section2FFound=false;
    }
    else
    {
      cout << "invalid section1F" << endl;
      pthread_exit (0);
    }
    }
    
    //validating section1F
    
    if(section1FPart1.length()>1 && section1FPart1[0]!=section1FPart2[1]){
        section1FPart2+=section1FPart1[0];
    }
    else{
      cout << "invalid section1F not long enough" << endl;
      pthread_exit (0);
    }
    
    for(int i=1; section1FPart1.length()>i;i++){
        //if last number equals current number cut out both numbers
        if(section1FPart1[i-1]==section1FPart1[i]){
            section1FPart2=section1FPart2.substr(0,section1FPart2.length()-1);
           break;
        }
        
        //if number has already been entered cut at the new number
        else if(section1FPart2.find(section1FPart1[i])!= string::npos){
           break;
        }
        else{
            section1FPart2+=section1FPart1[i];
        }
    }
     
    
    for(int i=0; section1FPart2.length()>i;i++){
        //if last number equals current number cut out both numbers
        if(section1FPart2[i]>n){
            n=section1FPart2[i];
        }
    }
       max= n;
     
      
      if(section1FPart2.find('0')!= string::npos){
            cout << "section1FPart2 contains a 0"<<endl;
           pthread_exit (0);
      }
      
      //check to make sure numbers from max to 1 are there 
   while(n>48){
       if(section1FPart2.find(n)!= string::npos){
       }
       else{
           cout << "section1FPart2 is not composed of numberical numbers from max to 1"<<endl;
           pthread_exit (0);
       }
      
       n--;
   }
   
   //convert from ascii value to integer for max
   max-=48;
     
     
        //validate section2F
 for (int j=0; j < section2F.length (); j++){
      
    if (isspace(section2F[j]) || isalpha(section2F[j])){
	    //read space
	}
    else {
      cout << "invalid section2F characters other than spaces or alpha " << endl;
      pthread_exit (0);
    }
    }
    //where j is the number of rows
    j=0;
 
    //check if section2F is divisible by the max
    if(section2F.length()%max==0){
        j=section2F.length()/max;
    }
    else{
        cout<< "invalid section2F length is not divisible by max"<<endl;
         pthread_exit (0);
    }
    
    char textMatr [j][max];
    int counter=0;
    
    //setup matrix 
    //j is the number of rows
    //max is the number of columns
    for(int l=0; l<max; l++){
        for(int p=0; p<j; p++){
            textMatr[p][l]=section2F[counter];
            counter++;
        }
    }
   
                //Swapping columns
                char textMatr_2 [j][max];
                for (int l = 0; l < max; l++)
                {
                    for (int p = 0; p<j; p++)
                    {
                        //take into account -1 -48 to convert from ascii -49
                        textMatr_2[p][l] =  textMatr[p][section1FPart2[l]-49];
                        
                    }
                }
               

        
 

  cout << "--------------Fence---------------" << endl;
         
               //print
                for (int l = 0; l < j; l++)
                {
                    for (int p = 0; p < max; p++)
                    {
                        cout << textMatr_2[l][p];
                    }
                }
                cout << endl;
                
  return EXIT_SUCCESS;
}

//Algorithm #2: Hill Algorithm
void *
hillT (void *in)
{
  string *input = (string *) (in);
  string message = *input;
  string encryptedMessage;
  bool section3HFound = false;
  int section1H;
  string section2H, section3H;

//read whitespace before section1H
  int i = 0;
  while (isspace (message[i]))
    {
      i++;
    }

  //get the message for section1H and checks b if the character in section1 is either 2 or 3
  if ((message[i] == '3' || message[i] == '2'))
    {

      //checks a if section1H is longer than one character
      if (isspace (message[i + 1]) || isalpha (message[i + 1]))
	{
	 
	}
      else
	{
	  cout << "invalid section1H is longer than one character" << endl;
	}
      section1H = message[i]-48;
    }
    else
    {
      cout << "invalid section1H does not equal 2 or 3." << endl;
      pthread_exit (0);
    }
    
    //increment once
    i++;
    
  //eliminate white space in front of section2H
  while (isspace (message[i]))
    {
      i++;
    }

  //get message 2 and 3 
  for (i; i < message.length () && section3HFound == false; i++)
    {
      if (isspace (message[i]))
      {
	
	  //read spaces
	}
      else if (isalpha (message[i]))
	{
	  section2H += message[i];
	}
      else if (isdigit (message[i]))
	{
	  section3H = message.substr(i, string::npos);
	  section3HFound = true;
	 
	}
      else
	{
	  //checks c if there is a non alphabet character in section2H
	  cout << "unexpected character in section2H" << endl;
	  pthread_exit (0);
	}
    }

  //convert to uppercase for section2H
  for (int j = 0; j < section2H.length (); j++)
    {
      section2H[j] = toupper (section2H[j]);

    }

  //validate sections

  //checks d
  if (section1H == 2 && section2H.length () % 2 != 0)
    {
      cout <<
	"section2H has wrong length. Should be a multiple of 2." << endl;
      pthread_exit (0);
    }
  if (section1H == 3 && section2H.length () % 3 != 0)
    {
      cout << "section2H has wrong length. Should be a multiple of 3." << endl;
      pthread_exit (0);
    }

  //checks e if section3H includes characters other than numerical characters
  for (int j = 0; j < section3H.length (); j++)
    {
      if (isdigit (section3H[j]) || (isspace (section3H[j])))
	{
	}
      else
	{

	  cout << "section3H contains non numberical characters." << endl;
	  pthread_exit (0);

	}
    }


  //Tokenize
  int *tokens = new int[section3H.length ()];
  char *cstr = new char[section3H.length () + 1];
  strcpy (cstr, section3H.c_str ());
  char *toks = strtok (cstr, " ");
  i = 0;
  while (toks != 0)
    {
      tokens[i] = atoi (toks);

      //cout << tokens[i] << " ";
      i++;
      toks = strtok (NULL, " ");
    }
  delete[]cstr;
  
  

  //checks f for more than 4 tokens if section1H is 2
  //encrypts
  if (section1H == 2 && i> 3)
    {
      for (int k = 0, m = 0; k < section2H.length (); k += 2, m++)
	{
	  encryptedMessage +=(char) (((((section2H[k]) - 65) * tokens[0] +(section2H[k + 1] - 65) * tokens[2]) % 26) + 65);
	  encryptedMessage += (char) (((((section2H[k]) - 65) * tokens[1] + (section2H[k + 1] - 65) * tokens[3]) % 26) + 65);
	}
    }
 
  //checks g for more than 9 tokens if section1H is 3
  else if (section1H == 3 && i >8)
    {
      for (int k = 0, m = 0; k < section2H.length (); k += 3, m++)
	{

	  encryptedMessage +=(char) (((((section2H[k]) - 65) * tokens[0] + (section2H[k + 1] - 65) * tokens[3] + (section2H[k + 2] -65) * tokens[6]) % 26) + 65);
	  encryptedMessage +=(char) (((((section2H[k]) - 65) * tokens[1] + (section2H[k + 1] - 65) * tokens[4] + (section2H[k +  2] - 65) * tokens[7]) % 26) + 65);
					  encryptedMessage +=(char) (((((section2H[k]) - 65) * tokens[2] +(section2H[k + 1] -65) * tokens[5] + (section2H[k + 2] - 65) * tokens[8]) % 26) + 65);

	}
    }
  else{
      if(section1H == 2){
          cout << "section3H does not have at least 4 tokens" <<endl;
          pthread_exit (0);
      }
      else if (section1H == 3){
          cout << "section3H does not have at least 9 tokens" << endl;
          pthread_exit (0);
      }
      else{
          cout << "error checking token length for hill section3H" <<endl;
          pthread_exit (0);
      }
  }
  
  cout << "--------------Hill----------------" << endl;
  cout << encryptedMessage << endl;

  pthread_exit (0);
  return EXIT_SUCCESS;
}

//Algorithm #3: Valley Algorithm
void *
valleyT (void *in)
{

  string *input = (string *) (in);
  string message = *input;
  string encryptedMessage;
  bool section3VFound = false;
  int section1V;
  string section2V, section3V;
  int determinant;
  int h;
  bool hFound=true;

//read whitespace before section1V
  int i = 0;
  while (isspace (message[i]))
    {
      i++;
    }

  //get the message for section1V and checks b if the character in section1V is either 2 or 3
  if ((message[i] == '3' || message[i] == '2'))
    {

      //checks a if section1V is longer than one character
      if (isspace(message[i + 1]) || isalpha (message[i + 1]))
	{
	 
	}
      else
	{
	  cout << "invalid section1V is longer than one character" << endl;
	}
      section1V = message[i]-48;
    }
    else
    {
      cout << "invalid section1V does not equal 2 or 3." << endl;
      pthread_exit (0);
    }
    
    //increment once
    i++;
    
  //eliminate white space in front of section2H
  while (isspace(message[i]))
    {
      i++;
    }

  //get message 2 and 3 
  for (i; i < message.length () && section3VFound == false; i++)
    {
      if (isspace(message[i]))
      {
	
	  //read spaces
	}
      else if (isalpha (message[i]))
	{
	  section2V += message[i];
	}
      else if (isdigit (message[i]))
	{
	  section3V = message.substr(i, string::npos);
	  section3VFound = true;
	 
	}
      else
	{
	  //checks c if there is a non alphabet character in section2V
	  cout << "unexpected character in section2V" << endl;
	  pthread_exit (0);
	}
    }

  //convert to uppercase for section2V
  for (int j = 0; j < section2V.length (); j++)
    {
      section2V[j] = toupper (section2V[j]);

    }

  //validate sections

  //checks d
  if (section1V == 2 && section2V.length () % 2 != 0)
    {
      cout <<
	"section2V has wrong length. Should be a multiple of 2." << endl;
      pthread_exit (0);
    }
  if (section1V == 3 && section2V.length () % 3 != 0)
    {
      cout << "section2 has wrong length. Should be a multiple of 3." << endl;
      pthread_exit (0);
    }

  //checks e if section3V includes characters other than numerical characters
  for (int j = 0; j < section3V.length (); j++)
    {
      if (isdigit (section3V[j]) || (isspace(section3V[j])))
	{
	}
      else
	{

	  cout << "section3V contains non numberical characters." << endl;
	  pthread_exit (0);

	}
    }


  //tokenize
  int *tokens = new int[section3V.length ()];
  int *tokensCopy = new int[section3V.length ()];
  char *cstr = new char[section3V.length () + 1];
  strcpy (cstr, section3V.c_str ());
  char *toks = strtok (cstr, " ");
  i = 0;
  while (toks != 0)
    { 
      tokens[i] = atoi (toks);
      tokensCopy[i] = atoi (toks);
      i++;
      toks = strtok (NULL, " ");
    }
  delete[]cstr;
  
  
  //encrypts
  
  
  //checks f for more than 4 tokens if section1V is 2
  if (section1V == 2 && i > 3)
    {
        //find the determinant of K
         determinant= tokens[0]*tokens[3]-tokens[1]*tokens[2];
         
          //find h 
      //for negative determinant add 26
      for (h=1; h<26; h++){
       if (determinant*h%26 +26 ==1){
            break;
       }
      }
      
      if(h==26){
          cout<<"Cannot compute inverse because section3V is not relatively prime."<< endl;
          pthread_exit(0);
      }
         
    
       //fill in each token using the formula
     tokens[0]=h*(tokensCopy[3]) % 26;
     tokens[1]=-h*(tokensCopy[1]) % 26;
     tokens[2]=-h*(tokensCopy[2]) % 26;
     tokens[3]=h*(tokensCopy[0]) % 26;
 
    
    //check if they are negative
    for (int k=0; k<section2V.length(); k++){
        if(tokens[k]<0){
            tokens[k]=tokens[k]+26;
        }
    }
     
      for (int k = 0, m = 0; k < section2V.length (); k += 2, m++)
	{
	  encryptedMessage +=(char) (((((section2V[k]) - 65) * tokens[0] + (section2V[k + 1] - 65) * tokens[2]) % 26) + 65);
	  encryptedMessage += (char) (((((section2V[k]) - 65) * tokens[1] +(section2V[k + 1] - 65) * tokens[3]) % 26) + 65);
	}
    }
    
    
  //checks g for more than 9 tokens if section1V is 3
  else if (section1V == 3 && i > 8)
    {
        //find the determinant of K
         determinant=tokens[0]*(tokens[4]*tokens[8]-tokens[5]*tokens[7])-tokens[1]*(tokens[3]*tokens[8]-tokens[5]*tokens[6])+tokens[2]*(tokens[3]*tokens[7]-tokens[4]*tokens[6]);
     
     
      //find h 
      //for negative determinant add 26
      for (h=1; h<26; h++){
       if (determinant*h%26 +26 ==1){
            break;
       }
      }
      
      if(h==26){
          cout<<"Cannot compute inverse because section3V is not relatively prime."<< endl;
          pthread_exit(0);
      }
   
      //fill in each token using the formula
     tokens[0]=h*(tokensCopy[4]*tokensCopy[8]-(tokensCopy[7]*tokensCopy[5])) % 26;
     tokens[1]=-h*(tokensCopy[1]*tokensCopy[8]-(tokensCopy[2]*tokensCopy[7])) % 26;
     tokens[2]=h*(tokensCopy[1]*tokensCopy[5]-tokensCopy[2]*tokensCopy[4]) % 26;
     tokens[3]=-h*(tokensCopy[3]*tokensCopy[8]-tokensCopy[5]*tokensCopy[6]) % 26;
     tokens[4]=h*(tokensCopy[0]*tokensCopy[8]-tokensCopy[2]*tokensCopy[6]) % 26;
     tokens[5]=-h*(tokensCopy[0]*tokensCopy[5]-tokensCopy[2]*tokensCopy[3]) % 26;
     tokens[6]=h*(tokensCopy[3]*tokensCopy[7]-tokensCopy[4]*tokensCopy[6]) % 26;
     tokens[7]=-h*(tokensCopy[0]*tokensCopy[7]-tokensCopy[1]*tokensCopy[6]) % 26;
     tokens[8]=h*(tokensCopy[0]*tokensCopy[4]-tokensCopy[1]*tokensCopy[3]) % 26;
    
    //check if they are negative
    for (int k=0; k<section2V.length(); k++){
        if(tokens[k]<0){
            tokens[k]=tokens[k]+26;
        }
    }
      
      //fill matrix with unencryptedMessage 
      for (int k = 0, m = 0; k < section2V.length (); k += 3, m++)
	{

	  encryptedMessage +=(char) (((((section2V[k]) - 65) * tokens[0] +(section2V[k + 1] -65) * tokens[3] + (section2V[k +2] -65) * tokens[6]) % 26) + 65);
	  encryptedMessage +=(char) (((((section2V[k]) - 65) * tokens[1] +(section2V[k + 1] -65) * tokens[4] + (section2V[k +2] -65) * tokens[7]) % 26) + 65);
	  encryptedMessage +=(char) (((((section2V[k]) - 65) * tokens[2] +(section2V[k + 1] -65) * tokens[5] + (section2V[k +2] -65) * tokens[8]) % 26) + 65);

	}
    }
else{
      if(section1V == 2){
          cout << "section3V does not have at least 4 tokens" <<endl;
          pthread_exit (0);
      }
      else if (section1V == 3){
          cout << "section3V does not have at least 9 tokens" <<endl;
          pthread_exit (0);
      }
      else{
          cout << "error checking token length for valley section3V"<<endl;
          pthread_exit (0);
      }
  }

  cout << "--------------Valley--------------" << endl;
  cout << encryptedMessage << endl;
  return EXIT_SUCCESS;
}
