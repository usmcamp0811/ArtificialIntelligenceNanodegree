"""Finish all TODO items in this file to complete the isolation project, then
test your agent's strength against a set of known agents using tournament.py
and include the results in your report.
"""
import random


class SearchTimeout(Exception):
    """Subclass base exception for code clarity. """
    pass

# check if p and o are on the same side of the vwall
def same_side_check(p_location, o_location, wall, axis):
    # 0 = left/up 1 = right/down
    p_orientation = []
    o_orientation = []
    for w in wall:
        if p_location[axis] < w:
            p_orientation.append(0)
        else:
            p_orientation.append(1)

        if o_location[axis] < w:
            o_orientation.append(0)
        else:
            o_orientation.append(1)
    return p_orientation == o_orientation

def custom_score(game, player):
    """Calculate the heuristic value of a game state from the point of view
    of the given player.

    This should be the best heuristic function for your project submission.

    Note: this function should be called from within a Player instance as
    `self.score()` -- you should not need to call this function directly.

    Parameters
    ----------
    game : `isolation.Board`
        An instance of `isolation.Board` encoding the current state of the
        game (e.g., player locations and blocked cells).

    player : object
        A player instance in the current game (i.e., an object corresponding to
        one of the player objects `game.__player_1__` or `game.__player_2__`.)

    Returns
    -------
    float
        The heuristic value of the current game state to the specified player.
    """

    # check if the game is over
    if game.is_winner(player):
        return float('inf')
    if game.is_loser(player):
        return float('-inf')

    n_player_moves = len(game.get_legal_moves(player))
    n_opponent_moves = len(game.get_legal_moves(game.get_opponent(player)))

    # make the agent more argressive as the game progresses
    percent_complete = len(game.get_blank_spaces()) / (game.height * game.width)

    aggression = 1
    if percent_complete <= 0.7:
        aggression += 0.15
    elif percent_complete <= .25:
        aggression += 0.25
    elif percent_complete <= .10:
        aggression += 0.5
    return n_player_moves - (aggression * n_opponent_moves)

def custom_score_2(game, player):
    """Calculate the heuristic value of a game state from the point of view
    of the given player.

    Note: this function should be called from within a Player instance as
    `self.score()` -- you should not need to call this function directly.

    Parameters
    ----------
    game : `isolation.Board`
        An instance of `isolation.Board` encoding the current state of the
        game (e.g., player locations and blocked cells).

    player : object
        A player instance in the current game (i.e., an object corresponding to
        one of the player objects `game.__player_1__` or `game.__player_2__`.)

    Returns
    -------
    float
        The heuristic value of the current game state to the specified player.
    """
    # check if the game is over
    if game.is_loser(player):
        return float("-inf")
    if game.is_winner(player):
        return float("inf")

    n_player_moves = len(game.get_legal_moves(player))
    n_opponent_moves = len(game.get_legal_moves(game.get_opponent(player)))

    # make the agent more argressive as the game progresses
    percent_complete = len(game.get_blank_spaces()) / (game.height * game.width)

    aggression = 1
    if percent_complete <= 0.7:
        aggression += 0.15
    elif percent_complete <= .25:
        aggression += 0.25
    elif percent_complete <= .10:
        aggression += 0.5

    # partition_score = board_partition_score(game, player)

    return (n_player_moves - n_opponent_moves)

def board_partition_score(game, player):
    """This method checks to see if the board has been partitioned and if
    both the players are on the same side of the partition or not. Made
    this originally without realizing the players are moving in an 'L' shape
    so it doesn't work as well as I would like.
    """
    # check if the game is over
    if game.is_loser(player):
        return float("-inf")
    if game.is_winner(player):
        return float("inf")

    # Check to see if the board is divided
    # if board is split:
    ## find out which side each player is on
        # if players on opposite sides:
        ##  count the number of moves... winner has most moves

    p_location = game.get_player_location(player)
    o_location = game.get_player_location(game.get_opponent(player))

    blanks = game.get_blank_spaces()
    rows, cols = zip(*blanks)

    rowscount = [rows.count(x) for x in range(game.height)]
    colscount = [cols.count(x) for x in range(game.width)]
    if 0 in colscount:
        vwall = [i for i, x in enumerate(colscount) if x == 0]
        vcheck = same_side_check(p_location, o_location, vwall, 1)
    else:
        vwall = False
        vcheck = True

    if 0 in rowscount:
        hwall = [i for i, x in enumerate(rowscount) if x == 0]
        hcheck = same_side_check(p_location, o_location, hwall, 0)
    else:
        hwall = False
        hcheck = True

    if vcheck & hcheck == True:
        return 1.0 # TODO: i dunno need to decided what is a good thing to return
    else:
        # Count the number of moves per player

        n_player_moves = len(game.get_legal_moves(player))
        n_opponent_moves = len(game.get_legal_moves(game.get_opponent(player)))
        if n_player_moves > n_opponent_moves:
            return n_player_moves
        else:
            return -n_player_moves

def custom_score_3(game, player):
    """Calculate the heuristic value of a game state from the point of view
    of the given player.

    Note: this function should be called from within a Player instance as
    `self.score()` -- you should not need to call this function directly.

    Parameters
    ----------
    game : `isolation.Board`
        An instance of `isolation.Board` encoding the current state of the
        game (e.g., player locations and blocked cells).

    player : object
        A player instance in the current game (i.e., an object corresponding to
        one of the player objects `game.__player_1__` or `game.__player_2__`.)

    Returns
    -------
    float
        The heuristic value of the current game state to the specified player.
    """
    if game.is_winner(player):
        return float('inf')
    if game.is_loser(player):
        return float('-inf')

    return float(len(game.get_legal_moves(player)))

class IsolationPlayer:
    """Base class for minimax and alphabeta agents -- this class is never
    constructed or tested directly.

    ********************  DO NOT MODIFY THIS CLASS  ********************

    Parameters
    ----------
    search_depth : int (optional)
        A strictly positive integer (i.e., 1, 2, 3,...) for the number of
        layers in the game tree to explore for fixed-depth search. (i.e., a
        depth of one (1) would only explore the immediate sucessors of the
        current state.)

    score_fn : callable (optional)
        A function to use for heuristic evaluation of game states.

    timeout : float (optional)
        Time remaining (in milliseconds) when search is aborted. Should be a
        positive value large enough to allow the function to return before the
        timer expires.
    """
    def __init__(self, search_depth=9, score_fn=custom_score, timeout=10.):
        self.search_depth = search_depth
        self.score = score_fn
        self.time_left = None
        self.TIMER_THRESHOLD = timeout


class MinimaxPlayer(IsolationPlayer):
    """Game-playing agent that chooses a move using depth-limited minimax
    search. You must finish and test this player to make sure it properly uses
    minimax to return a good move before the search time limit expires.
    """

    def get_move(self, game, time_left):
        """Search for the best move from the available legal moves and return a
        result before the time limit expires.

        **************  YOU DO NOT NEED TO MODIFY THIS FUNCTION  *************

        For fixed-depth search, this function simply wraps the call to the
        minimax method, but this method provides a common interface for all
        Isolation agents, and you will replace it in the AlphaBetaPlayer with
        iterative deepening search.

        Parameters
        ----------
        game : `isolation.Board`
            An instance of `isolation.Board` encoding the current state of the
            game (e.g., player locations and blocked cells).

        time_left : callable
            A function that returns the number of milliseconds left in the
            current turn. Returning with any less than 0 ms remaining forfeits
            the game.

        Returns
        -------
        (int, int)
            Board coordinates corresponding to a legal move; may return
            (-1, -1) if there are no available legal moves.
        """
        self.time_left = time_left

        # Initialize the best move so that this function returns something
        # in case the search fails due to timeout
        best_move = (-1, -1)

        try:
            # The try/except block will automatically catch the exception
            # raised when the timer is about to expire.
            return self.minimax(game, self.search_depth)

        except SearchTimeout:
            return best_move

        # Return the best move from the last completed search iteration
        return best_move

    def minimax(self, game, depth):
        """Implement depth-limited minimax search algorithm as described in
        the lectures.

        This should be a modified version of MINIMAX-DECISION in the AIMA text.
        https://github.com/aimacode/aima-pseudocode/blob/master/md/Minimax-Decision.md

        **********************************************************************
            You MAY add additional methods to this class, or define helper
                 functions to implement the required functionality.
        **********************************************************************

        Parameters
        ----------
        game : isolation.Board
            An instance of the Isolation game `Board` class representing the
            current game state

        depth : int
            Depth is an integer representing the maximum number of plies to
            search in the game tree before aborting

        Returns
        -------
        (int, int)
            The board coordinates of the best move found in the current search;
            (-1, -1) if there are no legal moves

        Notes
        -----
            (1) You MUST use the `self.score()` method for board evaluation
                to pass the project tests; you cannot call any other evaluation
                function directly.

            (2) If you use any helper functions (e.g., as shown in the AIMA
                pseudocode) then you must copy the timer check into the top of
                each helper function or else your agent will timeout during
                testing.
        """
        if self.time_left() < self.TIMER_THRESHOLD:
            raise SearchTimeout()

        # get possible moves
        player_moves = game.get_legal_moves()
        best_move = (-1,-1)
        # check to see if moves remain
        if not player_moves:
            # print('no moves')
            return (-1,-1)


        best_score = float('-inf')


        # step through and evaluate all possible moves at this level
        for current_move in player_moves:
            # print(current_move)
            forecast_game = game.forecast_move(current_move)
            # go deeper...
            score = max(best_score, self.min_value(forecast_game, depth))
            if score > best_score:
                best_score = score
                best_move = current_move

        return best_move

    def min_value(self, game, depth):

        if self.time_left() < self.TIMER_THRESHOLD:
            raise SearchTimeout()

        if depth == 0:
            return self.score(game, self)
        player_moves = game.get_legal_moves()
        best_score = float('inf')
        for move in player_moves:
            forecast_game = game.forecast_move(move)
            best_score = max(best_score, self.max_value(forecast_game, depth-1))

        return best_score

    def max_value(self, game, depth):

        if self.time_left() < self.TIMER_THRESHOLD:
            raise SearchTimeout()

        if depth == 0:
            return self.score(game, self)

        player_moves = game.get_legal_moves()
        best_score = float('-inf')

        for move in player_moves:
            forecast_game = game.forecast_move(move)
            best_score = max(best_score, self.min_value(forecast_game, depth-1))

        return best_score


class AlphaBetaPlayer(IsolationPlayer):
    """Game-playing agent that chooses a move using iterative deepening minimax
    search with alpha-beta pruning. You must finish and test this player to
    make sure it returns a good move before the search time limit expires.
    """

    def get_move(self, game, time_left):
        """Search for the best move from the available legal moves and return a
        result before the time limit expires.

        Modify the get_move() method from the MinimaxPlayer class to implement
        iterative deepening search instead of fixed-depth search.

        **********************************************************************
        NOTE: If time_left() < 0 when this function returns, the agent will
              forfeit the game due to timeout. You must return _before_ the
              timer reaches 0.
        **********************************************************************

        Parameters
        ----------
        game : `isolation.Board`
            An instance of `isolation.Board` encoding the current state of the
            game (e.g., player locations and blocked cells).

        time_left : callable
            A function that returns the number of milliseconds left in the
            current turn. Returning with any less than 0 ms remaining forfeits
            the game.

        Returns
        -------
        (int, int)
            Board coordinates corresponding to a legal move; may return
            (-1, -1) if there are no available legal moves.
        """

        self.time_left = time_left
        # have a move ready
        best_move = (-1, -1)

        try:
            depth = 1
            # while time_left() < self.TIMER_THRESHOLD:
                # print('time: ', time_left)
            best_move = self.alphabeta(game, depth)
            depth += 1
            # print('no timeout: ', best_move)
            return best_move

        except SearchTimeout:
            # print('timeout: ', best_move)
            return best_move

    def alphabeta(self, game, depth, alpha=float("-inf"), beta=float("inf")):
        """Implement depth-limited minimax search with alpha-beta pruning as
        described in the lectures.

        This should be a modified version of ALPHA-BETA-SEARCH in the AIMA text
        https://github.com/aimacode/aima-pseudocode/blob/master/md/Alpha-Beta-Search.md

        **********************************************************************
            You MAY add additional methods to this class, or define helper
                 functions to implement the required functionality.
        **********************************************************************

        Parameters
        ----------
        game : isolation.Board
            An instance of the Isolation game `Board` class representing the
            current game state

        depth : int
            Depth is an integer representing the maximum number of plies to
            search in the game tree before aborting

        alpha : float
            Alpha limits the lower bound of search on minimizing layers

        beta : float
            Beta limits the upper bound of search on maximizing layers

        Returns
        -------
        (int, int)
            The board coordinates of the best move found in the current search;
            (-1, -1) if there are no legal moves

        Notes
        -----
            (1) You MUST use the `self.score()` method for board evaluation
                to pass the project tests; you cannot call any other evaluation
                function directly.

            (2) If you use any helper functions (e.g., as shown in the AIMA
                pseudocode) then you must copy the timer check into the top of
                each helper function or else your agent will timeout during
                testing.
        """

        if self.time_left() < self.TIMER_THRESHOLD:
            raise SearchTimeout()

        # get possible moves
        player_moves = game.get_legal_moves()
        best_move = (-1, -1)
        # check to see if moves remain
        if not player_moves:
            # print('no moves')
            return (-1, -1)

        best_score = float('-inf')

        # step through and evaluate all possible moves at this level
        for current_move in player_moves:
            # print(current_move)
            forecast_game = game.forecast_move(current_move)
            # print('go deeper...')
            score = max(best_score, self.min_value(forecast_game, depth, alpha, beta))
            if score > best_score:
                best_score = score
                best_move = current_move

        alpha = max(alpha, best_score)
        if best_score >= beta:
            return best_move

        return best_move

    def min_value(self, game, depth, alpha, beta):

        if self.time_left() < self.TIMER_THRESHOLD:
            raise SearchTimeout()

        if depth == 0:
            return self.score(game, self)

        player_moves = game.get_legal_moves()
        best_score = float('inf')

        for move in player_moves:
            forecast_game = game.forecast_move(move)
            score = self.max_value(forecast_game, depth - 1, alpha, beta)
            if score < best_score:
                best_score = score

            if best_score <= alpha:
                return best_score

            # update beta
            beta = min(beta, best_score)

        return best_score

    def max_value(self, game, depth, alpha, beta):

        if self.time_left() < self.TIMER_THRESHOLD:
            raise SearchTimeout()

        if depth == 0:
            return self.score(game, self)

        player_moves = game.get_legal_moves()
        best_score = float('-inf')

        for move in player_moves:
            forecast_game = game.forecast_move(move)
            score = self.min_value(forecast_game, depth - 1, alpha, beta)
            if score > best_score:
                best_score = score

            if best_score >= alpha:
                return best_score

            # update alpha
            alpha = max(alpha, best_score)

        return best_score
