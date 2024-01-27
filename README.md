# project-1
the first project
void read_proposed_combination(enum color_t board[]) {
  for (int i = 0; i < NUM_PAWNS; i++) {
    char color_input[15];
    printf("Enter color for pawn %d (RED, GREEN, BLUE, YELLOW, BLACK, WHITE, GRAY, PURPLE): ", i + 1);
    scanf("%s", color_input);

    if (strcmp(color_input, "RED") == 0) {
      board[i] = RED;
    } else if (strcmp(color_input, "GREEN") == 0) {
      board[i] = GREEN;
    } // ... continue for other colors
    else {
      printf("Invalid color, try again.\n");
      i--;  
    }
  }
}


void evaluate_proposed_combination(
    enum color_t hidden_combination[],
    enum color_t proposed_combination[],
    int *num_well_placed_pawns,
    int *num_misplaced_pawns) {
  *num_well_placed_pawns = 0;
  *num_misplaced_pawns = 0;

  for (int i = 0; i < NUM_PAWNS; i++) {
    if (hidden_combination[i] == proposed_combination[i]) {
      (*num_well_placed_pawns)++;
    } else {

      for (int j = 0; j < NUM_PAWNS; j++) {
        if (hidden_combination[i] == proposed_combination[j]) {
          (*num_misplaced_pawns)++;
          break;
        }
      }
    }
  }
}

void game() {
  enum color_t hidden_combination[NUM_PAWNS];
  enum color_t proposed_combination[NUM_PAWNS];


  generate_hidden_combination(hidden_combination);

  int attempt = 1;
  while (attempt <= NUM_ATTEMPTS) {
    printf("\nAttempt %d:\n", attempt);


    read_proposed_combination(proposed_combination);

    int num_well_placed_pawns, num_misplaced_pawns;
    evaluate_proposed_combination(hidden_combination, proposed_combination, &num_well_placed_pawns, &num_misplaced_pawns);


    printf("%d well-placed pawns, %d misplaced pawns.\n", num_well_placed_pawns, num_misplaced_pawns);

    if (num_well_placed_pawns == NUM_PAWNS) {
      printf("Congratulations! You guessed the code!\n");
      break;
    }

    attempt++;
  }

  if (attempt > NUM_ATTEMPTS) {
    printf("You ran out of attempts. The hidden combination was:\n");

  }
}
