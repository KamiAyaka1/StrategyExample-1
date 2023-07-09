# Simple Strategy Game

# Set up the game board
board = [
  ['-', '-', '-'],
  ['-', '-', '-'],
  ['-', '-', '-']
]

# Define the players
players = ['X', 'O']
current_player = players[0]

# Game loop
loop do
  # Display the current game board
  puts "\nCurrent Board:"
  board.each { |row| puts row.join(' ') }

  # Prompt the current player for their move
  puts "\n#{current_player}'s turn. Enter the row and column numbers (e.g., '1 2'):"
  move = gets.chomp.split.map(&:to_i)

  # Validate the move
  if move.length != 2 || !move.all? { |m| m.between?(0, 2) } || board[move[0]][move[1]] != '-'
    puts "Invalid move. Please try again."
    next
  end

  # Update the game board with the player's move
  board[move[0]][move[1]] = current_player

  # Check for a winning condition
  winning_combinations = [
    [[0, 0], [0, 1], [0, 2]], # Row 1
    [[1, 0], [1, 1], [1, 2]], # Row 2
    [[2, 0], [2, 1], [2, 2]], # Row 3
    [[0, 0], [1, 0], [2, 0]], # Column 1
    [[0, 1], [1, 1], [2, 1]], # Column 2
    [[0, 2], [1, 2], [2, 2]], # Column 3
    [[0, 0], [1, 1], [2, 2]], # Diagonal top-left to bottom-right
    [[0, 2], [1, 1], [2, 0]]  # Diagonal top-right to bottom-left
  ]

  if winning_combinations.any? { |combo| combo.all? { |pos| board[pos[0]][pos[1]] == current_player } }
    puts "\n#{current_player} wins!"
    break
  end

  # Check for a tie condition
  if board.flatten.none? { |pos| pos == '-' }
    puts "\nIt's a tie!"
    break
  end

  # Switch to the next player
  current_player = (current_player == players[0]) ? players[1] : players[0]
end
