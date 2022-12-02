# AI_Dynamic_Environment
## Setup
Once you have cloned the repository onto your machine, simply move or copy the `environment.py` file into the directory where your jupyter notebook is being run. From there, you can simply add `from environment import truly_dynamic_environment, replay` to gain access to these two functions.
## Assumptions that I made about the agents
I made a couple of assumptions about the agents that would be playing together in order for this to work smoothly. The first is that your agent accepts the current board state as the first argument formatted as a 2d numpy array. The second assumption that I made about the agents is that they return a python dictionary where one of the keys is "move" and the value for that key is an integer value representing the column that the agent wishes to place a piece in (0 indexed). For example, my agents return a dictionary that looks something like this(as long as the move key exists, this environment should work):
```asm
{
    "move":5,
    "value":0.563
}
```
## Usage
### Truly_dynamic_environment
The function header for `truly_dynamic_environment` is as follows:
`truly_dynamic_environment(players,size=(6,7),visual=False,board=None)`
Where the players is a python dictionary containing information about the algorithms that will be playing against each other. Size is the board size, defaulted to 6x7. Visual is a boolean attribute that allows you to view the agents play eachother in realtime. ___note: the visual attribute will only work in jupyter notebook, not a regular python file.___ And board is the board to start from, setting this value to None will generate a fresh board for the agents to play on.
#### Players dictionary
The players dictionary should be formatted as follows:
```asm
players = [
    {
        "algo":<player_1_agent_function>,
        "player":1,
        "args":{<a dictionary of the other arguments to pass to the function>}
    },
    {
        "algo":<player_2_agent_function>,
        "player":-1,
        "args":{<a dictionary of the other arguments to pass to the function>}
    },
    
]
```
For example, to play my alpha beta pruning agent against my monte carlo agent, I would create players as follows:
```asm
players = [
    {
        "algo":a_b_cutoff_search,
        "player":1,
        "args":{
            "verbose":False,
            "cutoff":7,
            "player":1,
            "eval_func":monte_carlo_threaded_eval
        }
    },
    {
        "algo":pmcs,
        "player":-1,
        "args":{
            "N":3600,
            "verbose":False,
            "player":-1,
            "playout_func":random_player
        }
    },
    
]
```
Where the function headers for my alpha beta cutoff and pure monte carlo look like `a_b_cutoff_search(board,cutoff=None,player=1,verbose=False,eval_func=HelperFunctions.evaluate_board)` and `pmcs(board,N=50,player=1,verbose=False,playout_func=random_player)` respectively. 

#### Return types
The dynamic environment function returns 3 objects. The first is a python dictionary formatted as follows:
```asm
result = {
    "algo_info": {
        "<Algo_1_name>: {
            "time": <array of the time it took the algo to move per turn>
        }
    },
    "turns_taken":<total number of turns taken by both agents>,
    "winner": <1 or -1 depending on which algo won the game>
} 
```
The second is the final board where one of the agents won and the third is a list containing all board states that the agents got into.
### Replay
This function simply allows the user to view the game played by the agents and accepts a list of all board states and optionally, a sleep value. The sleep value is how long the funciton waits before showing you the next move that an agent made and is defaulted to 1. __note that, similar to the visual attribute in the environment, this function is designed to only work in Jupyter Notebook__


