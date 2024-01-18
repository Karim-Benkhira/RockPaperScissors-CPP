# Rock-Paper-Scissors Game

This C++ program implements a Rock-Paper-Scissors game where the player competes against the computer. The game allows multiple rounds, displays the choices made by both players, and announces the winner of each round. At the end of the game, overall statistics are presented.

## Program Structure

### Enums
- `enGamechoice`: Represents the choices in the game (Stone, Paper, Scissors).
- `enWinner`: Represents the possible winners of a round (Player1, Computer, Draw).

### Structures
- `stRoundInfo`: Holds information about each round, including the round number, choices made by both players, round winner, and winner's name.
- `stGameResults`: Stores the overall game results, including the number of rounds played, wins for both players, draw times, the overall game winner, and the winner's name.

### Functions
- `RandomNumber(int From, int To)`: Generates a random number within a specified range.
- `WinnerName(enWinner Winner)`: Converts the winner enum to a string representation.
- `WhoWonTheRound(stRoundInfo RoundInfo)`: Determines the winner of a round based on player choices.
- `choiceName(enGamechoice Choice)`: Converts the choice enum to a string representation.
- `SetWinnerScreenColor(enWinner Winner)`: Sets console color based on the winner.
- `PrintRoundResults(stRoundInfo RoundInfo)`: Prints the results of each round.
- `WhoWonTheGame(short Player1WinTimes, short ComputerWintimes)`: Determines the overall game winner.
- `FillGameResults(int GameRounds, short Player1WinTimes, short ComputerWintimes, short Drawtimes)`: Fills the `stGameResults` structure.
- `ReadPlayer1Choice()`: Reads the player's choice from the console.
- `GetComputerChoice()`: Generates a random choice for the computer.
- `PlayGame(short HowManyRounds)`: Manages the main gameplay loop, updating game statistics.
- `Tabs(short NumberofTabs)`: Creates a string with tabs for formatting.
- `ShowGameOverScreen()`: Displays the game-over screen.
- `ShowFinalGameResults(stGameResults GameResults)`: Prints the final game results.
- `ReadHowManyRounds()`: Reads the number of rounds to play from the user.
- `ResetScreen()`: Clears the console screen.
- `startGame()`: Initiates the game loop.

### Main
- Initializes the random seed.
- Calls the `startGame()` function to begin the game loop.

## How to Play

1. Ensure you have a C++ compiler installed on your system.
2. Clone the repository to your local machine.
3. Compile the program using your preferred C++ compiler.
4. Run the executable.

## Rock-Paper-Scissors Game

```cpp
#include <iostream>
#include <cstdlib>

using namespace std;

enum enGamechoice {Stone = 1,Paper = 2,Scissors = 3};
enum enWinner {Player1 = 1,Computer = 2,Draw = 3};

struct stRoundInfo
{
    short RoundNumber = 0;
    enGamechoice Player1choice;
    enGamechoice ComputerChoice;
    enWinner Winner;
    string WinnerName;
};

struct stGameResults
{
    short GameRounds = 0;
    short Player1WinTimes = 0;
    short Computer2Wintimes = 0;
    short Drawtimes = 0;
    enWinner GameWinner;
    string WinnerName = "";
};

int RandomNumber(int From,int To)
{
    int RandNB = rand() % (To - From + 1) + From;
    return RandNB;
}

string WinnerName(enWinner Winner)
{

    string arrwinnerName[3] = {"Player1","Computer","No winner"};
    return arrwinnerName[Winner - 1];
}

enWinner WhoWonTheRound(stRoundInfo RoundInfo)
{
    if(RoundInfo.Player1choice == RoundInfo.ComputerChoice)
    {
        return enWinner::Draw;
    }

    switch (RoundInfo.Player1choice)
    {
    case enGamechoice::Stone:
        if(RoundInfo.ComputerChoice == enGamechoice::Paper)
        {
            return enWinner::Computer;
        }
        break;
    case enGamechoice::Paper:
        if(RoundInfo.ComputerChoice == enGamechoice::Scissors)
        {
            return enWinner::Computer;
        }
        break;
    case enGamechoice::Scissors:
        if(RoundInfo.ComputerChoice == enGamechoice::Stone)
        {
            return enWinner::Computer;
        }
        break;
    
    }
    return enWinner::Player1;
}

string choiceName(enGamechoice Choice)
{
    string arrGameChoice[3] = {"Stone","Paper","Scissors"};
    return arrGameChoice[Choice-1];
}

void SetWinnerScreenColor(enWinner Winner)
{
    switch ((Winner))
    {
    case enWinner::Player1:
        system("color 2F");
        break;
    case enWinner::Computer:
        system("color 4F");
        cout << "\a";
        break;
    
    default:
        system("color 6F");
        break;
    }
}

void PrintRoundResults(stRoundInfo RoundInfo)
{
    cout << "\n____________Round [" << RoundInfo.RoundNumber << "]____________\n\n";
    cout << "Player1 Choice: " << choiceName(RoundInfo.Player1choice) << endl;
    cout << "Coputer Choice: " << choiceName(RoundInfo.ComputerChoice) << endl;
    cout << "Round Winner   : [" << RoundInfo.WinnerName << "]\n";
    cout << "__________________________________\n" << endl;
    SetWinnerScreenColor(RoundInfo.Winner);

}

enWinner WhoWonTheGame(short Player1WinTimes,short computerWintimes)
{
    if(Player1WinTimes > computerWintimes)
        return enWinner::Player1;
    else if(computerWintimes > Player1WinTimes)
        return enWinner::Computer;
    else
        return enWinner::Draw;
}

stGameResults FillGameResults(int GameRounds,short Player1WinTimes,short computerWintimes,short Drawtimes)
{
    stGameResults GameResults;

    GameResults.GameRounds = GameRounds;
    GameResults.Player1WinTimes = Player1WinTimes;
    GameResults.Computer2Wintimes = computerWintimes;
    GameResults.Drawtimes = Drawtimes;
    GameResults.GameWinner = WhoWonTheGame(Player1WinTimes,computerWintimes);
    GameResults.WinnerName = WinnerName(GameResults.GameWinner);

    return GameResults;
}

enGamechoice ReadPlayer1Choice()
{
    short Choice = 1;

    do
    {
        cout << "\nYour Choice: [1]:Stone, [2]:Paper, [3]:Scissors ? ";
        cin >> Choice;
    } while (Choice < 1 || Choice > 3);

    return (enGamechoice)Choice;
    
}

enGamechoice GetComputerChoice()
{
    return (enGamechoice) RandomNumber(1,3);
}

stGameResults PlayGame(short HowManyRonds)
{
    stRoundInfo RoundInfo;
    short Player1WinTimes = 0,ComputerWintimes = 0, DrawTimes = 0;

    for(short GameRound = 1;GameRound <= HowManyRonds;GameRound++)
    {
        cout << "\nRound [" << GameRound << "] begins:\n";
        RoundInfo.RoundNumber = GameRound;
        RoundInfo.Player1choice = ReadPlayer1Choice();
        RoundInfo.ComputerChoice = GetComputerChoice();
        RoundInfo.Winner = WhoWonTheRound(RoundInfo);
        RoundInfo.WinnerName = WinnerName(RoundInfo.Winner);

        if(RoundInfo.Winner == enWinner::Player1)
            Player1WinTimes++;
        else if (RoundInfo.Winner == enWinner::Computer)
            ComputerWintimes++;
        else
            DrawTimes++;

        PrintRoundResults(RoundInfo);
    }
    return FillGameResults(HowManyRonds,Player1WinTimes,ComputerWintimes,DrawTimes);
}

string Tabs(short NumberofTabs)
{
    string t = "";

    for(int i= 0;i < NumberofTabs;i++)
    {
        t = t + "\t";
        cout << t;
    }
    return t;
}

void ShowGameOverScreen()
{
    cout << Tabs(2) << "__________________________________________________________\n\n";
    cout << Tabs(2) << "                 +++ G a m e  O v e r +++\n";
    cout << Tabs(2) << "__________________________________________________________\n\n";
}

void ShowFinalGameResults(stGameResults GameResults)
{
    cout << Tabs(2) << "_____________________ [Game Results ]_____________________\n\n";
    cout << Tabs(2) << "Game Rounds        : " << GameResults.GameRounds << endl;
    cout << Tabs(2) << "Player1 won times  : " << GameResults.Player1WinTimes << endl;
    cout << Tabs(2) << "Computer won times : " << GameResults.Computer2Wintimes << endl;
    cout << Tabs(2) << "Draw times         : " << GameResults.Drawtimes << endl;
    cout << Tabs(2) << "Final Winner       : " << GameResults.WinnerName << endl;
    cout << Tabs(2) << "___________________________________________________________\n"; 
    SetWinnerScreenColor(GameResults.GameWinner);
}

short ReadHowManyRounds()
{
    short GameRounds = 1;

    do
    {
        cout << "How Many Rounds 1 to 10 ? \n";
        cin >> GameRounds;
    } while (GameRounds < 1 || GameRounds > 10);

    return GameRounds;
}

void ResetScreen()
{
    system("cls");
    system("color 0F");
}

void startGame()
{
    char PlayAgain = 'Y';

    do
    {
        ResetScreen();
        stGameResults GameResults = PlayGame(ReadHowManyRounds());
        ShowGameOverScreen();
        ShowFinalGameResults(GameResults);

        cout << endl << Tabs(3) << "Do you want to play again? Y/N? ";

        cin >> PlayAgain;
    } while (PlayAgain == 'Y' || PlayAgain == 'y');
    
}

int main()
{
    srand((unsigned)time(NULL));

    startGame();

    return 0;
}

```

### Author

- **Name:** Karim Benkhira
- **Occupation:** Software Engineering Student
- **LinkedIn:** [karim-benkhira](https://linkedin.com/in/karim-benkhira-206597224)
- **GitHub:** [Karim-Benkhira](https://github.com/Karim-Benkhira)
- **Twitter:** [Karim_Benkhira](https://twitter.com/Karim_Benkhira)

### Cybersecurity Enthusiast

I am also passionate about ethical hacking and cybersecurity. Currently, I am a beginner in penetration testing and constantly learning new techniques to enhance my skills.